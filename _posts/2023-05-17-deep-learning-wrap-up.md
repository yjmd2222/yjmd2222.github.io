---
title:  "AIB 딥러닝 섹션 회고"
excerpt: "딥러닝 제대로 배운 것도 없는데 끝남"
header:
  # teaser:
categories: [AIB_Section_3]
tags: [AIB, 딥러닝, deep_learning]
date: 2023-05-17 20:00
last_modified_at: 2023-05-17T20:00:00-09:00
---

한 달 동안 딥러닝을 배웠지만, 너무 방대한 양이므로 정리하기도 힘들다. 내가 얻은 교훈은...마음을 잘 정리해야 한다. 몇 년 걸려서 배울 내용을 한 달 안에 억지로 욱여 넣으면 당연히 안 되는데 그걸 진행하고 있는 부트캠프들은 대단하다. 왜 하나의 딥러닝 세부분야를 골라서 면밀히 가르치지 않고 "이런 것이 있습니다"라면서 매일 같이 새로운 것을 던져 주는지 이해할 수 없다. 마음을 비우고 시간 날 때 하나씩 여러 예제를 찾아보는 것이 좋다. 노트도 어차피 TensorFlow 튜토리얼 그대로 가져오는데, 최신 버전도 아니고 예전 버전이다. 계속 에러 난다. "노답" 이라는 생각을 저 구석에 치워 놓고 스스로 배워야 한다. 별로 좋지 않은 교육자료로 스스로 좋은 자료를 찾아보도록 자기주도학습을 유도한 것인가 싶다.

마음 한 쪽 답답했던 문제가 하나 있었는데 오늘 아침에 해결했다. 그 문제는 N332의 U-Net에서 *왜 몇 개의 layer만 골라서 모델로 구성하는지*였다. [튜토리얼](https://www.tensorflow.org/tutorials/images/segmentation?hl=ko)
```python
base_model = tf.keras.applications.MobileNetV2(input_shape=[128, 128, 3], include_top=False)

# Use the activations of these layers
layer_names = [
    'block_1_expand_relu',   # 64x64
    'block_3_expand_relu',   # 32x32
    'block_6_expand_relu',   # 16x16
    'block_13_expand_relu',  # 8x8
    'block_16_project',      # 4x4
]
base_model_outputs = [base_model.get_layer(name).output for name in layer_names]

# Create the feature extraction model
down_stack = tf.keras.Model(inputs=base_model.input, outputs=base_model_outputs)

down_stack.trainable = False
```
딱 이 부분인데, `layer_names`로 다섯 개의 layer만 구성한 인코더로 보인다. 사전학습 모델에서 중간 중간 사용할 layer만 선택하는 것으로 보이는 문제. 그런데 `down_stack.summary()`를 보면 마지막 layer까지 다 들어 있다.

```python
                                                                     
 block_3_pad (ZeroPadding2D)    (None, 33, 33, 144)  0           ['block_3_expand_relu[0][0]']    
                                                                                                  
 block_3_depthwise (DepthwiseCo  (None, 16, 16, 144)  1296       ['block_3_pad[0][0]']            
 nv2D)                                                                                            
                                                                                                  
 block_3_depthwise_BN (BatchNor  (None, 16, 16, 144)  576        ['block_3_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_3_depthwise_relu (ReLU)  (None, 16, 16, 144)  0           ['block_3_depthwise_BN[0][0]']   
                                                                                                  
 block_3_project (Conv2D)       (None, 16, 16, 32)   4608        ['block_3_depthwise_relu[0][0]'] 
                                                                                                  
 block_3_project_BN (BatchNorma  (None, 16, 16, 32)  128         ['block_3_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_4_expand (Conv2D)        (None, 16, 16, 192)  6144        ['block_3_project_BN[0][0]']     
                                                                                                  
 block_4_expand_BN (BatchNormal  (None, 16, 16, 192)  768        ['block_4_expand[0][0]']         
 ization)                                                                                         
                                                                                                  
 block_4_expand_relu (ReLU)     (None, 16, 16, 192)  0           ['block_4_expand_BN[0][0]']      
                                                                                                  
 block_4_depthwise (DepthwiseCo  (None, 16, 16, 192)  1728       ['block_4_expand_relu[0][0]']    
 nv2D)                                                                                            
                                                                                                  
 block_4_depthwise_BN (BatchNor  (None, 16, 16, 192)  768        ['block_4_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_4_depthwise_relu (ReLU)  (None, 16, 16, 192)  0           ['block_4_depthwise_BN[0][0]']   
                                                                                                  
 block_4_project (Conv2D)       (None, 16, 16, 32)   6144        ['block_4_depthwise_relu[0][0]'] 
                                                                                                  
 block_4_project_BN (BatchNorma  (None, 16, 16, 32)  128         ['block_4_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_4_add (Add)              (None, 16, 16, 32)   0           ['block_3_project_BN[0][0]',     
                                                                  'block_4_project_BN[0][0]']     
                                                                                                  
 block_5_expand (Conv2D)        (None, 16, 16, 192)  6144        ['block_4_add[0][0]']            
                                                                                                  
 block_5_expand_BN (BatchNormal  (None, 16, 16, 192)  768        ['block_5_expand[0][0]']         
 ization)                                                                                         
                                                                                                  
 block_5_expand_relu (ReLU)     (None, 16, 16, 192)  0           ['block_5_expand_BN[0][0]']      
                                                                                                  
 block_5_depthwise (DepthwiseCo  (None, 16, 16, 192)  1728       ['block_5_expand_relu[0][0]']    
 nv2D)                                                                                            
                                                                                                  
 block_5_depthwise_BN (BatchNor  (None, 16, 16, 192)  768        ['block_5_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_5_depthwise_relu (ReLU)  (None, 16, 16, 192)  0           ['block_5_depthwise_BN[0][0]']   
                                                                                                  
 block_5_project (Conv2D)       (None, 16, 16, 32)   6144        ['block_5_depthwise_relu[0][0]'] 
                                                                                                  
 block_5_project_BN (BatchNorma  (None, 16, 16, 32)  128         ['block_5_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_5_add (Add)              (None, 16, 16, 32)   0           ['block_4_add[0][0]',            
                                                                  'block_5_project_BN[0][0]']     
                                                                                                  
 block_6_expand (Conv2D)        (None, 16, 16, 192)  6144        ['block_5_add[0][0]']            
                                                                                                  
 block_6_expand_BN (BatchNormal  (None, 16, 16, 192)  768        ['block_6_expand[0][0]']         
 ization)                                                                                         
                                                                                                  
 block_6_expand_relu (ReLU)     (None, 16, 16, 192)  0           ['block_6_expand_BN[0][0]']      
                                                                                                  
 block_6_pad (ZeroPadding2D)    (None, 17, 17, 192)  0           ['block_6_expand_relu[0][0]']    
                                                                                                  
 block_6_depthwise (DepthwiseCo  (None, 8, 8, 192)   1728        ['block_6_pad[0][0]']            
 nv2D)                                                                                            
                                                                                                  
 block_6_depthwise_BN (BatchNor  (None, 8, 8, 192)   768         ['block_6_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_6_depthwise_relu (ReLU)  (None, 8, 8, 192)    0           ['block_6_depthwise_BN[0][0]']   
                                                                                                  
 block_6_project (Conv2D)       (None, 8, 8, 64)     12288       ['block_6_depthwise_relu[0][0]'] 
                                                                                                  
 block_6_project_BN (BatchNorma  (None, 8, 8, 64)    256         ['block_6_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_7_expand (Conv2D)        (None, 8, 8, 384)    24576       ['block_6_project_BN[0][0]']     
                                                                                                  
 block_7_expand_BN (BatchNormal  (None, 8, 8, 384)   1536        ['block_7_expand[0][0]']         
 ization)                                                                                         
                                                                                                  
 block_7_expand_relu (ReLU)     (None, 8, 8, 384)    0           ['block_7_expand_BN[0][0]']      
                                                                                                  
 block_7_depthwise (DepthwiseCo  (None, 8, 8, 384)   3456        ['block_7_expand_relu[0][0]']    
 nv2D)                                                                                            
                                                                                                  
 block_7_depthwise_BN (BatchNor  (None, 8, 8, 384)   1536        ['block_7_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_7_depthwise_relu (ReLU)  (None, 8, 8, 384)    0           ['block_7_depthwise_BN[0][0]']   
                                                                                                  
 block_7_project (Conv2D)       (None, 8, 8, 64)     24576       ['block_7_depthwise_relu[0][0]'] 
                                                                                                  
 block_7_project_BN (BatchNorma  (None, 8, 8, 64)    256         ['block_7_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_7_add (Add)              (None, 8, 8, 64)     0           ['block_6_project_BN[0][0]',     
                                                                  'block_7_project_BN[0][0]']     
                                                                                                  
 block_8_expand (Conv2D)        (None, 8, 8, 384)    24576       ['block_7_add[0][0]']            
                                                                                                  
 block_8_expand_BN (BatchNormal  (None, 8, 8, 384)   1536        ['block_8_expand[0][0]']         
 ization)                                                                                         
                                                                                                  
 block_8_expand_relu (ReLU)     (None, 8, 8, 384)    0           ['block_8_expand_BN[0][0]']      
                                                                                                  
 block_8_depthwise (DepthwiseCo  (None, 8, 8, 384)   3456        ['block_8_expand_relu[0][0]']    
 nv2D)                                                                                            
                                                                                                  
 block_8_depthwise_BN (BatchNor  (None, 8, 8, 384)   1536        ['block_8_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_8_depthwise_relu (ReLU)  (None, 8, 8, 384)    0           ['block_8_depthwise_BN[0][0]']   
                                                                                                  
 block_8_project (Conv2D)       (None, 8, 8, 64)     24576       ['block_8_depthwise_relu[0][0]'] 
                                                                                                  
 block_8_project_BN (BatchNorma  (None, 8, 8, 64)    256         ['block_8_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_8_add (Add)              (None, 8, 8, 64)     0           ['block_7_add[0][0]',            
                                                                  'block_8_project_BN[0][0]']     
                                                                                                  
 block_9_expand (Conv2D)        (None, 8, 8, 384)    24576       ['block_8_add[0][0]']            
                                                                                                  
 block_9_expand_BN (BatchNormal  (None, 8, 8, 384)   1536        ['block_9_expand[0][0]']         
 ization)                                                                                         
                                                                                                  
 block_9_expand_relu (ReLU)     (None, 8, 8, 384)    0           ['block_9_expand_BN[0][0]']      
                                                                                                  
 block_9_depthwise (DepthwiseCo  (None, 8, 8, 384)   3456        ['block_9_expand_relu[0][0]']    
 nv2D)                                                                                            
                                                                                                  
 block_9_depthwise_BN (BatchNor  (None, 8, 8, 384)   1536        ['block_9_depthwise[0][0]']      
 malization)                                                                                      
                                                                                                  
 block_9_depthwise_relu (ReLU)  (None, 8, 8, 384)    0           ['block_9_depthwise_BN[0][0]']   
                                                                                                  
 block_9_project (Conv2D)       (None, 8, 8, 64)     24576       ['block_9_depthwise_relu[0][0]'] 
                                                                                                  
 block_9_project_BN (BatchNorma  (None, 8, 8, 64)    256         ['block_9_project[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_9_add (Add)              (None, 8, 8, 64)     0           ['block_8_add[0][0]',            
                                                                  'block_9_project_BN[0][0]']     
                                                                                                  
 block_10_expand (Conv2D)       (None, 8, 8, 384)    24576       ['block_9_add[0][0]']            
                                                                                                  
 block_10_expand_BN (BatchNorma  (None, 8, 8, 384)   1536        ['block_10_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_10_expand_relu (ReLU)    (None, 8, 8, 384)    0           ['block_10_expand_BN[0][0]']     
                                                                                                  
 block_10_depthwise (DepthwiseC  (None, 8, 8, 384)   3456        ['block_10_expand_relu[0][0]']   
 onv2D)                                                                                           
                                                                                                  
 block_10_depthwise_BN (BatchNo  (None, 8, 8, 384)   1536        ['block_10_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_10_depthwise_relu (ReLU)  (None, 8, 8, 384)   0           ['block_10_depthwise_BN[0][0]']  
                                                                                                  
 block_10_project (Conv2D)      (None, 8, 8, 96)     36864       ['block_10_depthwise_relu[0][0]']
                                                                                                  
 block_10_project_BN (BatchNorm  (None, 8, 8, 96)    384         ['block_10_project[0][0]']       
 alization)                                                                                       
                                                                                                  
 block_11_expand (Conv2D)       (None, 8, 8, 576)    55296       ['block_10_project_BN[0][0]']    
                                                                                                  
 block_11_expand_BN (BatchNorma  (None, 8, 8, 576)   2304        ['block_11_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_11_expand_relu (ReLU)    (None, 8, 8, 576)    0           ['block_11_expand_BN[0][0]']     
                                                                                                  
 block_11_depthwise (DepthwiseC  (None, 8, 8, 576)   5184        ['block_11_expand_relu[0][0]']   
 onv2D)                                                                                           
                                                                                                  
 block_11_depthwise_BN (BatchNo  (None, 8, 8, 576)   2304        ['block_11_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_11_depthwise_relu (ReLU)  (None, 8, 8, 576)   0           ['block_11_depthwise_BN[0][0]']  
                                                                                                  
 block_11_project (Conv2D)      (None, 8, 8, 96)     55296       ['block_11_depthwise_relu[0][0]']
                                                                                                  
 block_11_project_BN (BatchNorm  (None, 8, 8, 96)    384         ['block_11_project[0][0]']       
 alization)                                                                                       
                                                                                                  
 block_11_add (Add)             (None, 8, 8, 96)     0           ['block_10_project_BN[0][0]',    
                                                                  'block_11_project_BN[0][0]']    
                                                                                                  
 block_12_expand (Conv2D)       (None, 8, 8, 576)    55296       ['block_11_add[0][0]']           
                                                                                                  
 block_12_expand_BN (BatchNorma  (None, 8, 8, 576)   2304        ['block_12_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_12_expand_relu (ReLU)    (None, 8, 8, 576)    0           ['block_12_expand_BN[0][0]']     
                                                                                                  
 block_12_depthwise (DepthwiseC  (None, 8, 8, 576)   5184        ['block_12_expand_relu[0][0]']   
 onv2D)                                                                                           
                                                                                                  
 block_12_depthwise_BN (BatchNo  (None, 8, 8, 576)   2304        ['block_12_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_12_depthwise_relu (ReLU)  (None, 8, 8, 576)   0           ['block_12_depthwise_BN[0][0]']  
                                                                                                  
 block_12_project (Conv2D)      (None, 8, 8, 96)     55296       ['block_12_depthwise_relu[0][0]']
                                                                                                  
 block_12_project_BN (BatchNorm  (None, 8, 8, 96)    384         ['block_12_project[0][0]']       
 alization)                                                                                       
                                                                                                  
 block_12_add (Add)             (None, 8, 8, 96)     0           ['block_11_add[0][0]',           
                                                                  'block_12_project_BN[0][0]']    
                                                                                                  
 block_13_expand (Conv2D)       (None, 8, 8, 576)    55296       ['block_12_add[0][0]']           
                                                                                                  
 block_13_expand_BN (BatchNorma  (None, 8, 8, 576)   2304        ['block_13_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_13_expand_relu (ReLU)    (None, 8, 8, 576)    0           ['block_13_expand_BN[0][0]']     
                                                                                                  
 block_13_pad (ZeroPadding2D)   (None, 9, 9, 576)    0           ['block_13_expand_relu[0][0]']   
                                                                                                  
 block_13_depthwise (DepthwiseC  (None, 4, 4, 576)   5184        ['block_13_pad[0][0]']           
 onv2D)                                                                                           
                                                                                                  
 block_13_depthwise_BN (BatchNo  (None, 4, 4, 576)   2304        ['block_13_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_13_depthwise_relu (ReLU)  (None, 4, 4, 576)   0           ['block_13_depthwise_BN[0][0]']  
                                                                                                  
 block_13_project (Conv2D)      (None, 4, 4, 160)    92160       ['block_13_depthwise_relu[0][0]']
                                                                                                  
 block_13_project_BN (BatchNorm  (None, 4, 4, 160)   640         ['block_13_project[0][0]']       
 alization)                                                                                       
                                                                                                  
 block_14_expand (Conv2D)       (None, 4, 4, 960)    153600      ['block_13_project_BN[0][0]']    
                                                                                                  
 block_14_expand_BN (BatchNorma  (None, 4, 4, 960)   3840        ['block_14_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_14_expand_relu (ReLU)    (None, 4, 4, 960)    0           ['block_14_expand_BN[0][0]']     
                                                                                                  
 block_14_depthwise (DepthwiseC  (None, 4, 4, 960)   8640        ['block_14_expand_relu[0][0]']   
 onv2D)                                                                                           
                                                                                                  
 block_14_depthwise_BN (BatchNo  (None, 4, 4, 960)   3840        ['block_14_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_14_depthwise_relu (ReLU)  (None, 4, 4, 960)   0           ['block_14_depthwise_BN[0][0]']  
                                                                                                  
 block_14_project (Conv2D)      (None, 4, 4, 160)    153600      ['block_14_depthwise_relu[0][0]']
                                                                                                  
 block_14_project_BN (BatchNorm  (None, 4, 4, 160)   640         ['block_14_project[0][0]']       
 alization)                                                                                       
                                                                                                  
 block_14_add (Add)             (None, 4, 4, 160)    0           ['block_13_project_BN[0][0]',    
                                                                  'block_14_project_BN[0][0]']    
                                                                                                  
 block_15_expand (Conv2D)       (None, 4, 4, 960)    153600      ['block_14_add[0][0]']           
                                                                                                  
 block_15_expand_BN (BatchNorma  (None, 4, 4, 960)   3840        ['block_15_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_15_expand_relu (ReLU)    (None, 4, 4, 960)    0           ['block_15_expand_BN[0][0]']     
                                                                                                  
 block_15_depthwise (DepthwiseC  (None, 4, 4, 960)   8640        ['block_15_expand_relu[0][0]']   
 onv2D)                                                                                           
                                                                                                  
 block_15_depthwise_BN (BatchNo  (None, 4, 4, 960)   3840        ['block_15_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_15_depthwise_relu (ReLU)  (None, 4, 4, 960)   0           ['block_15_depthwise_BN[0][0]']  
                                                                                                  
 block_15_project (Conv2D)      (None, 4, 4, 160)    153600      ['block_15_depthwise_relu[0][0]']
                                                                                                  
 block_15_project_BN (BatchNorm  (None, 4, 4, 160)   640         ['block_15_project[0][0]']       
 alization)                                                                                       
                                                                                                  
 block_15_add (Add)             (None, 4, 4, 160)    0           ['block_14_add[0][0]',           
                                                                  'block_15_project_BN[0][0]']    
                                                                                                  
 block_16_expand (Conv2D)       (None, 4, 4, 960)    153600      ['block_15_add[0][0]']           
                                                                                                  
 block_16_expand_BN (BatchNorma  (None, 4, 4, 960)   3840        ['block_16_expand[0][0]']        
 lization)                                                                                        
                                                                                                  
 block_16_expand_relu (ReLU)    (None, 4, 4, 960)    0           ['block_16_expand_BN[0][0]']     
                                                                                                  
 block_16_depthwise (DepthwiseC  (None, 4, 4, 960)   8640        ['block_16_expand_relu[0][0]']   
 onv2D)                                                                                           
                                                                                                  
 block_16_depthwise_BN (BatchNo  (None, 4, 4, 960)   3840        ['block_16_depthwise[0][0]']     
 rmalization)                                                                                     
                                                                                                  
 block_16_depthwise_relu (ReLU)  (None, 4, 4, 960)   0           ['block_16_depthwise_BN[0][0]']  
                                                                                                  
 block_16_project (Conv2D)      (None, 4, 4, 320)    307200      ['block_16_depthwise_relu[0][0]']
                                                                                                  
==================================================================================================
Total params: 1,841,984
Trainable params: 0
Non-trainable params: 1,841,984
__________________________________________________________________________________________________
```

`down_stack`으로 모델 저장할 때 `outputs=base_model_outputs`로 하면 `down_stack`의 `outputs`은 다섯 개의 layer의 output이 출력되고, 이것은 `down_stack.layers`의 각각의 output과는 다르다. 이렇게 5 layer의 outputs을 정의했으니, 쉽게 가져다 사용할 수 있게 된다:
```python
def unet_model(output_channels:int):
  inputs = tf.keras.layers.Input(shape=[128, 128, 3])

  # Downsampling through the model
  skips = down_stack(inputs)
  x = skips[-1]
  skips = reversed(skips[:-1])

  # Upsampling and establishing the skip connections
  for up, skip in zip(up_stack, skips):
    x = up(x)
    concat = tf.keras.layers.Concatenate()
    x = concat([x, skip])

  # This is the last layer of the model
  last = tf.keras.layers.Conv2DTranspose(
      filters=output_channels, kernel_size=3, strides=2,
      padding='same')  #64x64 -> 128x128

  x = last(x)

  return tf.keras.Model(inputs=inputs, outputs=x)
```

기타 모델 구성하는 것도 어느 정도 익숙해졌다. 왜 이렇게 구성해야 하는지 모르겠지만, layer 넣으라면 넣을 수 있을 정도. 섹션 3 프로젝트에서 autoencoder 잠깐 시도해봤는데, 모델 중간 레이어를 뽑아서 그걸 loss로 넣어보는 것도 해봤다. 다른 것도 여러 번 해보면 잘 되겠지.

이번 한 달간 몸이 너무 안 좋은데 때려치고 싶은 생각도 많이 들었다. 왜 이제까지 컴퓨터 앞에 앉아도 자세 관리 잘 했었는데 뭐가 달라졌는지 모르겠다. 허리 아픈것부터 시작해서 뒷목이 너무 땡기니 뭘 할 수가 없다. 의자에 기대어 앉으면 내 몸이 스스로 근육에 힘을 주어 앉는 것이 아니라서 일부터 딱딱하고 등도 기댈 수 없는 의자에 앉는데 비싼 돈주고 컴퓨터 의자 사야할 것 같다. 몸이 조금 괜찮았다가 다시 나빠졌다가 하는데 정말 스트레스다.