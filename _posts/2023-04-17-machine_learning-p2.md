---
title:  "AIB Section 2 머신러닝 Part 2"
excerpt: "지난 한 달 동안 배운 머신러닝 라이브러리"
header:
  teaser: /assets/images/s2ml00-machine-learning.png
categories:
  - AIB_Section_2
tags:
  - AIB
  - 머신러닝
  - machine_learning
date: 2023-04-17 15:00
last_modified_at: 2023-04-17T15:00:00-09:00
---

<p align="center">
  <img src="/assets/images/s2ml00-machine-learning.png" />
</p>

[Section 2 머신러닝 Part 1](/{% for subcategory in page.categories %}{{subcategory}}{% endfor %}/machine-learning-p1)에서 Section 2에서 배운 개념 내용을 정리했다. 이 글에서는 사용한 함수들을 목록으로 나열해보겠다. 지금은 목록으로만 작성하지만 다음 번에는 매일 배운 내용 정리해야겠다.

```python
# regression 알고리즘
from sklearn.linear_model import LinearRegression, Ridge, Lasso, ElasticNet, LogisticRegression
# 회귀문제 타겟 형태 변형
from sklearn.compose import TransformedTargetRegressor
# pipeline 안에 preprocessing과 모델 입력
from sklearn.pipeline import make_pipeline
# degree parameter로 받는 preprocessing. pipeline으로 LinearRegression과 연결 
from sklearn.preprocessing import PolynomialFeatures
# 평가지표 라이브러리
from sklearn.metrics import *
# 훈련, 테스트셋으로 나눌 수 있는 함수.
from sklearn.model_selection import train_test_split
# fold로 나누어 CV 진행. cross_val_score가 코드가 더 짧음.
from sklearn.model_selection import KFold
# Cross-validation
from sklearn.model_selection import cross_val_score
# 규제화 전 스케일링
from sklearn.preprocessing import StandardScaler, MinMaxScaler, KBinsDiscretizer, QuantileTransformer
# 결측치 대체
from sklearn.impute import SimpleImputer
# ordinal, one-hot encoder. category_encoders는 cols를 지정해주면 지정된 cols만 encoding됨. 코랩에서 별도 설치 필요
from category_encoders import OrdinalEncoder, OneHotEncoder, CountEncoder, TargetEncoder
# 점수 높은 feature 선택. 점수 함수 입력 필요
from sklearn.feature_selection import SelectKBest
# 칼럼에 대해 상세하게 설명: 결측치, 분포 등
from pandas_profiling import ProfileReport
# Decision Tree
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor
# tree plot
from sklearn.tree import export_graphviz
# Random Forest
from sklearn.ensemble import RandomForestClassifier, RandomForestRegressor
# XGB 별도 설치 필요
from xgboost import XGBClassifier, XGBRegressor
# LGBM 별도 설치 필요. XGB보다 훨씬 빠름
from lightgbm import LGBMClassifier, LGBMRegressor
# 하이퍼파라미터 튜닝
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV
# 별도 설치 Bayesian. 비교적 최신이라고 하긴 해도 GridSearchCV 성능 못 따라옴. 별도 함수 직접 작성해야 함
from hyperopt import hp
# 상관관계 확인 중 스피어만: 선형성만 아닌 단조성 확인 가능
from scipy.stats import spearmanr
# PI 별도 설치
from eli5.sklearn import PermutationImportance
# pdp
from pdpbox.pdp import pdp_isolate, pdp_plot, pdp_interact, pdp_interact_plot
# sampling 기법. 별도 설치 필요
from imblearn.under_sampling import RandomUnderSampler, RandomOverSampler, SMOTE, SMOTEENN
# ensemble sampling 접목. 위와 마찬가지로 같이 설치.
from imblearn.ensemble import BalancedBaggingClassifier
# 멀티클래스. 기본 알고리즘이 멀티클래스가 가능하지만 평가지표 자세히 살피기 위해 변형 필요
from sklearn.multiclass import OneVsRestClassifier
# 모델 저장
import pickle
```