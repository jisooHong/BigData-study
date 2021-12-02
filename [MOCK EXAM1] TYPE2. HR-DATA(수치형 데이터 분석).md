## [MOCK EXAM1] TYPE2. HR-DATA 
수치형 데이터만 분석하기 ( 찐입문자용 )   
-> object형 분석하기 힘들 때는 이거라도 하자

```python3
import pandas as pd
import numpy as np

X_test = pd.read_csv("../input/hr-data/X_test.csv")
X_train = pd.read_csv("../input/hr-data/X_train.csv")
y_train = pd.read_csv("../input/hr-data/y_train.csv")

# 각 필드의 데이터 타입 확인하기
print(X_train.info())

# 수치형 데이터만 선택하기
train = X_train.select_types(exclude = 'object')
test = X_test.select_types(excluse = 'object')

# 훈련되지 않아도 될 필드 삭제
train = train.drop('enrollee_id',axis=1)
test = test.drop('enrollee_id',axis=1)

# Training
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier()

model.fit(train,y_train['target'])
pred = model.predict_proba(test)[:,1] # proba 하면 이직하지 않을 확률(0일 확률), 이직 할 확률(1일 확률) 나옴 , 이중 1번째 값 선택

# 결과제출
pd.DataFrame({'enrollee_id': X_test.enrollee_id, 'target': pred}).to_csv('003000000.csv', index=False)
```
