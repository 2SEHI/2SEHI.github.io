---
title:  "[Research Paper] Character Region Awareness for Text Detection(읽는중)"
excerpt: "naver clova 2019"
date: 2021-10-17
last_modified_at: 2021-10-17

use_math: true
comments: true

categories:
  - Research Paper
---
# Character Region Awareness for Text Detection



논문 링크 : [https://arxiv.org/abs/1904.01941](https://arxiv.org/abs/1904.01941)



## 0.Abstract

> Scene text detection methods based on neural networks have emerged recently and have shown promising results. Previous methods trained with rigid word-level bounding boxes exhibit limitations in representing the text region in an arbitrary shape. In this paper, we propose a new scene text detection method to effectively detect text area by exploring each character and affinity between characters. To overcome the lack of individual character level annotations, our proposed framework exploits both the given character-level annotations for synthetic images and the estimated character-level ground-truths for real images acquired by the learned interim model. In order to estimate affinity between characters, the network is trained with the newly proposed representation for affinity. Extensive experiments on six benchmarks, including the TotalText and CTW-1500 datasets which contain highly curved texts in natural images, demonstrate that our character-level text detection significantly outperforms the state-of-the-art detectors. According to the results, our proposed method guarantees high flexibility in detecting complicated scene text images, such as arbitrarily-oriented, curved, or deformed texts.

신경망을 기반으로 한 문자 탐지 방법론은 최근 등장하여 촉망받는 결과를 보여주고 있다. 까다로운 문자 단위의 바운딩 박스로 훈련된 이전의 방법론은 독단적인 형태에서 문자영역을 대표하는 한계를 보여준다. 본 논문에서는, 각 문자의 탐색과 문자사이의 밀접한 관계를 통한 효율적인 새로운 문자탐지 방법론을 제안한다. 독립적인 문자 단위로 annotation 된 데이터 부족을 극복하기 위해서 합성이미지에 대해 주어진 문자 단위의 annotation과 학습중인 모델로 추정한 실제 이미지의 문자 단위 ground-truths을  활용한다. 문자간의 밀접한 관계를 추론하기 위해서 네트워크는 새롭게 제안된 밀접관계 표현으로 훈련되었다. 6개의 기준으로 행해진 광범위한 실험은 전체 문자와 자연 이미지에서 휘어진 글자를 포함한 CTW-1500 데이터셋을 포함하며, 문자 단위의 텍스트 탐지는 상당히 뛰어난 최신 탐지기를 입증한다. 결과에 따르면, 원근왜곡, 곡선, 변형된 텍스트 같은 복잡한 텍스트 이미지도 매우 유연하게 탐지할 수 있다.

- ground-truths : 모델이 우리가 원하는 답으로 예측해주길 바라는 답
  - [https://mac-user-guide.tistory.com/m/14?category=882578](https://mac-user-guide.tistory.com/m/14?category=882578)



### 요약 

문자단위의 바운딩 박스로 훈련하던 이전 방식과 달리 문자와 문자 사이의 관계표현을 이용해 텍스트 탐지를 행했으며 이는 매우 뛰어난 성능을 보여준다



## 1. Introduction

> Scene text detection has attracted much attention in the computer vision field because of its numerous applications, such as instant translation, image retrieval, scene parsing, geo-location, and blind-navigation. Recently, scene text detectors based on deep learning have shown promising performance [8, 40, 21, 4, 11, 10, 12, 13, 17, 24, 25, 32, 26]. These methods mainly train their networks to localize word level bounding boxes. However, they may suffer in difficult cases, such as texts that are curved, deformed, or extremely long, which are hard to detect with a single bounding box. Alternatively, character-level awareness has many advantages when handling challenging texts by linking the successive characters in a bottom-up manner. Unfortunately,Figure 1. Visualization of character-level detection using CRAFT. (a) Heatmaps predicted by our proposed framework. (b) Detection results for texts of various shape. most of the existing text datasets do not provide character level annotations, and the work needed to obtain character level ground truths is too costly. In this paper, we propose a novel text detector that localizes the individual character regions and links the detected characters to a text instance. Our framework, referred to as CRAFT for Character Region Awareness For Text detection, is designed with a convolutional neural network producing the character region score and affinity score. The region score is used to localize individual characters in the image, and the affinity score is used to group each character into a single instance. To compensate for the lack of character-level annotations, we propose a weakly supervised learning framework that estimates character-level ground truths in existing real word-level datasets. Figure. 1 is a visualization of CRAFT’s results on various shaped texts. By exploiting character-level region awareness, texts in various shapes are easily represented. We demonstrate extensive experiments on ICDAR datasets [15, 14, 28] to validate our method, and the experiments show that the proposed method outperforms state-of-the-art text detectors. Furthermore, experiments on MSRATD500, CTW-1500, and Total Text datasets [36, 38, 3] show the high flexibility of the proposed method on complicated cases, such as long, curved, and/or arbitrarily shaped texts.

Scene text detection는 실시간 번역, 이미지 검색, 장면 구문 분석, 지리 위치 와 블라인드 네비게이션 같이 많은 애플리케이션으로 인해 컴퓨터 비전 영역에서 관심을 받았다. 최근에 딥러닝을 기반으로 한 Scene text detectors 는 유망한 퍼포먼스를 보여준다. 

- 언급 논문
  - Single shot text detector with regional attention 
  - an efficient and accurate scene text detector
  - Fast oriented text spotting with a unified network
  - Detecting scene text via instance segmentation
  - Deep direct regression for multi-oriented scene text detection
  - Exploiting word annotations for character based text detection
  - An end-to-end textspotter with explicit alignment and attention
  - rotational region cnn for orientation robust scene text detection
  - A single-shot oriented scene text detector.
  - A flexible representation for detecting text of arbitrary shape
  - Mask textspotter: An end-to-end trainable neural network for spotting text with arbitrary shapes
  - Detecting oriented text in natural images by linking segments.
  - Multi-oriented scene text detection via corner localization and region segmentation







## 2. Discussions

> Robustness to Scale Variance We solely performed single scale experiments on all the datasets, even though the size of texts are highly diverse. This is different from the majority of other methods, which rely on multi-scale tests to handle the scale variance problem. This advantage comes from the property of our method localizing individual characters, not the whole text. The relatively small receptive field is sufficient to cover a single character in a large image, which makes CRAFT robust in detecting scale variant texts. Multi-language issue The IC17 dataset contains Bangla and Arabic characters, which are not included in the synthetic text dataset. Moreover, both languages are difficult to segment into characters individually because every character is written cursively. Therefore, our model could not distinguish Bangla and Arabic characters as well as it does Latin, Korean, Chinese, and Japanese. In East Asian characters’ cases, they can be easily separated with a constant width, which helps train the model to high performance via weakly-supervision. Comparison with End-to-end methods Our method is trained with the ground truth boxes only for detection, but it is comparable with other end-to-end methods, as shown in Table. 3. From the analysis of failure cases, we expect our model to benefit from the recognition results, especially when the ground truth words are separated by semantics, rather than visual cues. Generalization ability Our method achieved state-of-the art performances on 3 different datasets without additional fine-tuning. This demonstrates that our model is capable of capturing general characteristics of texts, rather than overfitting to a particular dataset.



## 3. Conclusion

> We have proposed a novel text detector called CRAFT, which can detect individual characters even when character level annotations are not given. The proposed method provides the character region score and the character affinity score that, together, fully cover various text shapes in a bottom-up manner. Since real datasets provided with character-level annotations are rare, we proposed a weakly supervised learning method that generates pseudo-ground truthes from an interim model. CRAFT shows state-of-the art performances on most public datasets and demonstrates generalization ability by showing these performances without fine-tuning. As our future work, we hope to train our model with a recognition model in an end-to-end fashion to see whether the performance, robustness, and generalizability of CRAFT translates to a better scene text spotting system that can be applied in more general settings. Acknowledgements. The authors would like to thank Beomyoung Kim, Daehyun Nam, and Donghyun Kim for helping with extensive experiments