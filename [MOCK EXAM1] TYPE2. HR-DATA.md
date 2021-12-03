[MOCK EXAM1] TYPE2. HR-DATA 최종
- 수치형 데이터 뿐만 아니라 범주형 데이터도 훈련시켜보자.
- 하이퍼 파라미터 튜닝으로 예측도를 향상시켜보자

< 주요 함수 >   
- ```select_dtypes(include = 'object').columns```   
```exclude``` 도 가능   
```columns``` 는 컬럼명을 반환   

```python3  
# 데이터 불러오기
import pandas as pd
import numpy as np

X_test = pd.read_csv("../input/hr-data/X_test.csv")
X_train = pd.read_csv("../input/hr-data/X_train.csv")
y_train = pd.read_csv("../input/hr-data/y_train.csv")


# 필요없는 데이터 삭제
train = X_train.drop('enrollee_id',axis=1)
test = X_test.drop('enrollee_id',axis=1)

# 범주형 데이터의 컬럼 이름 불러오기
colulmns = train.select_dtypes(include = 'object').columns

# 범주형 데이터 피처 엔지니어링
from sklearn.preprocessing import LabelEncoder
for col in columns:
  # NAN 처리
  train[col] = train[col].fillna('ignore')
  test[col] = test[col].fillna('ignore')
  
  le = LabelEncoder()
  train[col] = le.fit_transform(train[col])
  test[col] = le.fit_transform(test[col])

# Training
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators = 150, max_depth = 20, random_state = 2022)
model.fit(train,y_train['target'])

pred = model.predict_proba(test)[:,1]
pd.DataFrame({'enrollee_id': X_test.enrollee_id, 'target': pred}).to_csv('003000000.csv', index=False)
```

[TEST CODE]
```python3
# 체점(아래 주석 풀로 체점)
import pickle
import numpy as np
from sklearn.metrics import roc_auc_score

with open( "../input/hr-data/answer.pickle", "rb" ) as file:
    ans = pickle.load(file)
    ans = pd.DataFrame(ans)
    print(roc_auc_score(ans['target'], pred))
```
결과 : 0.7891509
